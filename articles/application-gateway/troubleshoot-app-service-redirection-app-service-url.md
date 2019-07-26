---
title: App Service ile Azure Application Gateway sorunlarını giderme – App Service URL 'sine yeniden yönlendirme
description: Bu makalede, Azure Application Gateway ile kullanıldığında yeniden yönlendirme sorununun nasıl giderileceği hakkında bilgi verilmektedir Azure App Service
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 07/19/2019
ms.author: absha
ms.openlocfilehash: 7baadfb549b19bb12757c82d723578202b5cf8ad
ms.sourcegitcommit: e72073911f7635cdae6b75066b0a88ce00b9053b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68347890"
---
# <a name="troubleshoot-app-service-issues-in-application-gateway"></a>Application Gateway App Service sorunlarını giderme

App Server Application Gateway ile arka uç hedefi olarak kullanıldığında karşılaşılan sorunları tanılamanıza ve çözmeyi öğrenin

## <a name="overview"></a>Genel Bakış

Bu makalede, aşağıdaki sorunları nasıl giderebileceğinizi öğreneceksiniz:

> [!div class="checklist"]
> * Yeniden yönlendirme olduğunda App Service URL 'SI tarayıcıda kullanıma alınıyor
> * App Service ARRAffinity tanımlama bilgisi etki alanı, özgün ana bilgisayar yerine App Service ana bilgisayar adı (example.azurewebsites.net) olarak ayarlandı

Bir arka uç uygulaması yeniden yönlendirme yanıtı gönderdiğinde, istemciyi arka uç uygulaması tarafından belirtilene göre farklı bir URL 'ye yönlendirmek isteyebilirsiniz. Örneğin, bir uygulama hizmeti bir uygulama ağ geçidinin arkasında barındırılıyorsa ve istemcinin göreli yoluna yeniden yönlendirme yapması için bunu yapmak isteyebilirsiniz. (Örneğin, contoso.azurewebsites.net/path1 öğesinden contoso.azurewebsites.net/path2 'e yeniden yönlendirme.) App Service bir yeniden yönlendirme yanıtı gönderdiğinde, uygulamanın konum üstbilgisindeki ana bilgisayar adını uygulama ağ geçidinden aldığı istekte olduğu gibi kullanır. Bu nedenle, istemci, uygulama ağ geçidi (contoso.com/path2) boyunca değil, isteği doğrudan contoso.azurewebsites.net/path2 'a yapar. Application Gateway 'i atlamak istenmez.

Bu sorun aşağıdaki başlıca nedenlerden kaynaklanıyor olabilir:

- App Service yeniden yönlendirme yapılandırdınız. Yeniden yönlendirme, isteğe bir sondaki eğik çizgi eklemek kadar basit olabilir.
- Yeniden yönlendirmeye neden olan Azure AD kimlik doğrulaması var.

Ayrıca, Application Gateway arka planda uygulama hizmetleri 'ni kullanırken, Application Gateway (example.com) ile ilişkili etki alanı adı App Service (deyin example.azurewebsites.net) etki alanı adından farklı olacaktır çünkü App Service tarafından ayarlanan ARRAffinity tanımlama bilgisinin etki alanı değeri, "example.azurewebsites.net" etki alanı adını taşır ve bu işlem istenmez. Özgün ana bilgisayar adı (example.com), tanımlama bilgisinde etki alanı adı değeri olmalıdır.

## <a name="sample-configuration"></a>Örnek yapılandırma

- HTTP dinleyicisi: Temel veya çok siteli
- Arka uç adres havuzu: App Service
- HTTP ayarları: "Arka uç adresinden ana bilgisayar adını seçin" etkin
- Yokla "HTTP ayarlarından konak adı Seç" etkin

## <a name="cause"></a>Nedeni

App Service çok kiracılı bir hizmet olduğundan, isteği doğru uç noktaya yönlendirmek için istekteki ana bilgisayar üst bilgisini kullanır. Ancak, uygulama hizmetlerinin varsayılan etki alanı adı *. azurewebsites.net (deyin contoso.azurewebsites.net), Application Gateway 'in etki alanı adından (deyin contoso.com) farklıdır. İstemciden gelen özgün istek ana bilgisayar adı olarak uygulama ağ geçidinin etki alanı adına (contoso.com) sahip olduğundan, uygulama ağ geçidini, isteği yönlendirdikleri sırada App Services 'in ana bilgisayar adına, özgün istekteki konak adını değiştirecek şekilde yapılandırmanız gerekir. App Service arka ucu.  Bunun için, Application Gateway HTTP ayarı yapılandırmasındaki "arka uç adresinden konak adresini seçin" anahtarını ve sistem durumu araştırması yapılandırmasındaki "arka uç HTTP ayarlarından ana bilgisayar adını seçin" anahtarını kullanarak elde edersiniz.



![appservice-1](./media/troubleshoot-app-service-redirection-app-service-url/appservice-1.png)

Bu nedenle, App Service bir yeniden yönlendirme yaparken, aksi belirtilmedikçe "contoso.com" özgün ana bilgisayar adı yerine "contoso.azurewebsites.net" geçersiz kılınan ana bilgisayar adını kullanır. Aşağıdaki örnek istek ve yanıt üst bilgilerini kontrol edebilirsiniz.
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
Yukarıdaki örnekte, Yanıt üstbilgisinin yeniden yönlendirme için 301 durum koduna sahip olduğunu ve konum üstbilgisinin özgün ana bilgisayar adı yerine App Service ana bilgisayar adının olduğunu fark edebilirsiniz.

## <a name="solution-rewrite-the-location-header"></a>Çözüm: Konum başlığını yeniden yazın

Konum üstbilgisindeki ana bilgisayar adını Application Gateway 'in etki alanı adına ayarlamanız gerekir. Bunu yapmak için, yanıttaki konum üstbilgisinin azurewebsites.net içerip içermediğini değerlendiren ve uygulama ağ geçidinin ana bilgisayar adına sahip olacak konum başlığını yeniden yazmak için bir eylem gerçekleştirerek bir [yeniden yazma kuralı](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers) oluşturun.  [Konum üst bilgisini yeniden yazma yönergeleri için](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers#modify-a-redirection-url)bkz.

> [!NOTE]
> HTTP üst bilgisi yeniden yazma desteği yalnızca Application Gateway [Standard_V2 ve WAF_v2 SKU 'su](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant) için kullanılabilir. V1 SKU 'SU kullanıyorsanız, v2 SKU 'SU ile kullanılabilen diğer [Gelişmiş özellikleri](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant#feature-comparison-between-v1-sku-and-v2-sku) kullanabilmek için [v1 'den v2 'ye geçiş](https://docs.microsoft.com/azure/application-gateway/migrate-v1-v2) yapmanızı önemle tavsiye ederiz.

## <a name="alternate-solution-use-app-services-custom-domain-instead-of-default-domain-name"></a>Alternatif çözüm: Varsayılan etki alanı adı yerine App Service 'in özel etki alanını kullan

V1 SKU kullanıyorsanız, bu yetenek yalnızca v2 SKU 'SU için kullanılabilir olduğundan konum başlığını yeniden yazmanız mümkün olmayacaktır. Bu nedenle, yeniden yönlendirme sorununu çözmek için Application Gateway App Service alınan aynı ana bilgisayar adını, bir konak geçersiz kılma işlemi yapmak yerine geçirmeniz gerekecektir.

Bunu yaptıktan sonra App Service, aynı orijinal ana bilgisayar üstbilgisinde yeniden yönlendirmeyi (varsa), Application Gateway işaret eder ve kendi kendine değil.

Bunu başarmak için bir özel etki alanına sahip olmanız ve aşağıda bahsedilen işlemi izlemeniz gerekir.

- Etki alanını App Service özel etki alanı listesine kaydedin. Bunun için, özel etki alanında App Service FQDN 'sine işaret eden bir CNAME olması gerekir. Daha fazla bilgi için bkz. [mevcut bir özel DNS adını Azure App Service eşleme](https://docs.microsoft.com//azure/app-service/app-service-web-tutorial-custom-domain).

![appservice-2](./media/troubleshoot-app-service-redirection-app-service-url/appservice-2.png)

- Bu işlem yapıldıktan sonra, App Service "www.contoso.com" ana bilgisayar adını kabul etmeye hazırlanın. Şimdi, DNS 'deki CNAME girdinizi Application Gateway FQDN 'sine geri işaret etmek üzere değiştirin. Örneğin, "appgw.eastus.cloudapp.azure.com".

- DNS sorgusu yaparken, "www.contoso.com" etki alanınız Application Gateway FQDN 'sine çözümlendiğinden emin olun.

- "Arka uç HTTP ayarlarından ana bilgisayar adını seçin" ayarını devre dışı bırakmak için özel araştırmayı ayarlayın Bu, set-AzApplicationGatewayProbeConfig komutunda-PickHostNameFromBackendHttpSettings anahtarı kullanılarak, araştırma ayarları ve PowerShell 'deki onay kutusunun işaretini kaldırarak portaldan yapılabilir. Araştırmanın konak adı alanında, Application Gateway gönderilen araştırma istekleri ana bilgisayar üstbilgisinde bunu taşıyacağından, App Service FQDN 'si "example.azurewebsites.net" değerini girin.

  > [!NOTE]
  > Sonraki adımı yaparken, lütfen bu noktada "arka uç adresinden ana bilgisayar seçin" anahtarı etkin olduğundan, özel araştırmanın arka uç HTTP ayarlarınızla ilişkili olmadığından emin olun.

- "Arka uç adresinden ana bilgisayar adını seçin" ayarını devre dışı bırakmak için Application Gateway HTTP ayarlarını ayarlayın. Bu, set-AzApplicationGatewayBackendHttpSettings komutunda-Pickhostnamefrombackendadddress anahtarı kullanılarak onay kutusu ve PowerShell 'in denetlenerek portaldan yapılabilir.

- Özel araştırmayı arka uç HTTP ayarlarına geri ilişkilendirin ve iyi durumda olduğunda arka uç durumunu doğrulayın.

- Bu işlem tamamlandıktan sonra, Application Gateway "www.contoso.com" ana bilgisayar adını App Service iletmesi gerekir ve yeniden yönlendirme aynı ana bilgisayar adı üzerinde gerçekleşecektir. Aşağıdaki örnek istek ve yanıt üst bilgilerini kontrol edebilirsiniz.

Mevcut bir kurulum için PowerShell 'i kullanarak yukarıda bahsedilen adımları uygulamak için aşağıdaki örnek PowerShell betiğini izleyin. Araştırma ve HTTP ayarları yapılandırmasındaki-PickHostname anahtarlarını nasıl kullandığımızda not edin.

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

Yukarıdaki adımlar sorunu çözmezse, bir [destek bileti](https://azure.microsoft.com/support/options/)açın.
