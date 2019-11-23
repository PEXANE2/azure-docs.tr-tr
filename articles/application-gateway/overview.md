---
title: Azure Application Gateway nedir?
description: Uygulamanıza web trafiğini yönetmek için bir Azure uygulama ağ geçidini nasıl kullanabileceğinizi öğrenin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: overview
ms.custom: mvc
ms.date: 11/23/2019
ms.author: victorh
ms.openlocfilehash: a61b1a44419ac35efa5888de2b5a6e4988dfb512
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74422305"
---
# <a name="what-is-azure-application-gateway"></a>Azure Application Gateway nedir?

Azure Application Gateway, web uygulamalarınıza trafiği yönetmenizi sağlayan bir web trafiği yük dengeleyicisidir. Geleneksel yük dengeleyiciler aktarım katmanında (OSI katman 4 - TCP ve UDP) çalışır ve trafiği kaynak IP adresi ve bağlantı noktasına göre hedef bir IP adresi ve bağlantı noktasına yönlendirir.

![Application Gateway conceptual](media/overview/figure1-720.png)

With Application Gateway, you can make routing decisions based on additional attributes of an HTTP request, such as URI path or host headers. Örneğin, gelen URL’yi temel alarak trafiği yönlendirebilirsiniz. Yani `/images` gelen URL’deyse, trafiği görüntüler için yapılandırılmış belirli bir sunucu kümesine (havuz olarak da bilinir) yönlendirebilirsiniz. If `/video` is in the URL, that traffic is routed to another pool that's optimized for videos.

![imageURLroute](./media/application-gateway-url-route-overview/figure1-720.png)

Bu yönlendirme türü, uygulama katmanı (OSI katman 7) yük dengelemesi olarak bilinir. Azure Application Gateway, URL tabanlı yönlendirmeyi ve daha fazlasını yapabilir.

>[!NOTE]
> Azure, senaryolarınız için tam olarak yönetilen yük dengeleme çözümleri sunar. If you need high-performance, low-latency, Layer-4 load balancing, see [What is Azure Load Balancer?](../load-balancer/load-balancer-overview.md) If you're looking for global DNS load balancing, see [What is Traffic Manager?](../traffic-manager/traffic-manager-overview.md) Your end-to-end scenarios may benefit from combining these solutions.
>
> For an Azure load-balancing options comparison, see [Overview of load-balancing options in Azure](https://docs.microsoft.com/azure/architecture/guide/technology-choices/load-balancing-overview).

Azure Application Gateway aşağıdaki özellikleri içerir:

## <a name="secure-sockets-layer-ssltls-termination"></a>Secure Sockets Layer (SSL/TLS) termination

Application gateway supports SSL/TLS termination at the gateway, after which traffic typically flows unencrypted to the backend servers. Bu özellik, web sunucularının maliyetli şifreleme ve şifre çözme ek yükünden kurtulmasını sağlar. But sometimes unencrypted communication to the servers is not an acceptable option. This can be because of security requirements, compliance requirements, or the application may only accept a secure connection. For these applications, application gateway supports end to end SSL/TLS encryption.

## <a name="autoscaling"></a>Otomatik ölçeklendirme

Application Gateway or WAF deployments under Standard_v2 or WAF_v2 SKU support autoscaling and can scale up or down based on changing traffic load patterns. Otomatik ölçeklendirme ayrıca sağlama sırasında dağıtım boyutu veya örnek sayısı seçme gereksinimini de ortadan kaldırır. For more information about the Application Gateway Standard_v2 and WAF_v2  features, see [Autoscaling v2 SKU](application-gateway-autoscaling-zone-redundant.md).

## <a name="zone-redundancy"></a>Zone redundancy

An Application Gateway or WAF deployments under  Standard_v2 or WAF_v2 SKU can span multiple Availability Zones, offering better fault resiliency and removing the need to provision separate Application Gateways in each zone.

## <a name="static-vip"></a>Static VIP

The application gateway VIP on Standard_v2 or WAF_v2 SKU supports static VIP type exclusively. This ensures that the VIP associated with application gateway doesn't change even over the lifetime of the Application Gateway.

## <a name="web-application-firewall"></a>Web uygulaması güvenlik duvarı

Web application firewall (WAF) is a service that provides centralized protection of your web applications from common exploits and vulnerabilities. WAF is based on rules from the [OWASP (Open Web Application Security Project) core rule sets](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) 3.1 (WAF_v2 only), 3.0, and 2.2.9. 

Web uygulamaları, bilinen yaygın güvenlik açıklarından yararlanan kötü amaçlı saldırıların giderek daha fazla hedefi olmaktadır. Bu açıklardan yararlanma örnekleri arasında SQL ekleme saldırıları, siteler arası komut dosyası saldırıları yaygındır. Uygulama kodunda bu tür saldırıların önlenmesi zor olabilir ve uygulama topolojisinin birçok katmanında ayrıntılı bakım, düzeltme eki uygulama ve izleme işlemleri gerektirebilir. Merkezi bir web uygulaması güvenlik duvarı, güvenlik yönetimini çok daha kolay hale getirir ve yetkisiz erişim ya da izinsiz giriş tehditlerine karşı uygulama yöneticilerine daha iyi güvence verir. Bir WAF çözümü, bilinen bir güvenlik açığına merkezi bir konumda düzeltme eki uygulayarak güvenlik tehdidine karşı, web uygulamalarının her birinin güvenliğini sağlamaya göre daha hızlı tepki verebilir. Var olan uygulama ağ geçitleri, web uygulaması güvenlik duvarı bulunan bir uygulama ağ geçidine kolaylıkla dönüştürülebilir.

For more information, see [What is Azure Web Application Firewall?](../web-application-firewall/overview.md).

## <a name="ingress-controller-for-aks"></a>AKS için Giriş Denetleyicisi
Application Gateway Ingress Controller (AGIC) allows you to use Application Gateway as the ingress for an [Azure Kubernetes Service (AKS)](https://azure.microsoft.com/services/kubernetes-service/) cluster. 

The ingress controller runs as a pod within the AKS cluster and consumes [Kubernetes Ingress Resources](https://kubernetes.io/docs/concepts/services-networking/ingress/) and converts them to an Application Gateway configuration which allows the gateway to load-balance traffic to the Kubernetes pods. The ingress controller only supports Application Gateway V2 SKU. 

For more information, see [Application Gateway Ingress Controller (AGIC)](ingress-controller-overview.md).

## <a name="url-based-routing"></a>URL tabanlı yönlendirme

URL Yolu Tabanlı Yönlendirme, trafiği isteğin URL Yollarına göre arka uç sunucu havuzlarına yönlendirmenizi sağlar. Senaryolardan biri, farklı içerik türleri için istekleri farklı havuzlara yönlendirmektir.

Örneğin, `http://contoso.com/video/*` için istekler VideoServerPool’a ve `http://contoso.com/images/*` için istekler ImageServerPool’a yönlendirilir. Yol desenlerinden hiçbiri eşleşmiyorsa DefaultServerPool seçilir.

For more information, see [URL-based routing with Application Gateway](https://docs.microsoft.com/azure/application-gateway/url-route-overview).

## <a name="multiple-site-hosting"></a>Birden çok site barındırma

Birden çok site barındırma, aynı uygulama ağ geçidi örneğinde birden fazla web sitesi yapılandırmanızı sağlar. This feature allows you to configure a more efficient topology for your deployments by adding up to 100 web sites to one Application Gateway, or 40 for WAF (for optimal performance). Her web sitesi kendi havuzuna yönlendirilebilir. Örneğin, uygulama ağ geçidi ContosoServerPool ve FabrikamServerPool adlı iki sunucu havuzundan `contoso.com` ve `fabrikam.com` için trafik sunabilir.

`http://contoso.com` için istekler ContosoServerPool’a ve `http://fabrikam.com` için istekler FabrikamServerPool’a yönlendirilir.

Benzer şekilde, aynı üst etki alanının iki alt etki alanı, aynı uygulama ağ geçidi dağıtımında barındırılabilir. Alt etki alanı kullanım örnekleri, tek bir uygulama ağ geçidi dağıtımında barındırılan `http://blog.contoso.com` ve `http://app.contoso.com` öğelerini içerebilir.

For more information, see [multiple-site hosting with Application Gateway](https://docs.microsoft.com/azure/application-gateway/multiple-site-overview).

## <a name="redirection"></a>Yönlendirme

Birçok web uygulaması için yaygın bir senaryo, bir uygulama ile kullanıcıları arasındaki tüm iletişimin şifrelenmiş bir yol üzerinden yapıldığından emin olmak için otomatik HTTP’yi HTTPS’ye dönüştürme yeniden yönlendirmesini desteklemektir.

In the past, you may have used techniques such as dedicated pool creation whose sole purpose is to redirect requests it receives on HTTP to HTTPS. Uygulama ağ geçidi, Application Gateway’de trafiği yeniden yönlendirme özelliğini destekler. Bu uygulama yapılandırmasını basitleştirir, kaynak kullanımını en iyi duruma getirir ve genel ve yol tabanlı yeniden yönlendirme dahil yeni yeniden yönlendirme senaryolarını destekler. Application Gateway redirection support isn't limited to HTTP to HTTPS redirection alone. Bu, kuralları kullanarak tanımladığınız herhangi bir bağlantı noktasından ve bağlantı noktasına yeniden yönlendirebilmeniz için genel bir yeniden yönlendirme mekanizmasıdır. Ayrıca, bir dış siteye yönlendirmeyi de destekler.

Application Gateway yeniden yönlendirme desteği aşağıdaki özellikleri sunar:

- Ağ Geçidi üzerindeki bir bağlantı noktasından başka bir bağlantı noktasına genel yeniden yönlendirme. Bu özellik, bir sitede HTTP’den HTTPS’ye yeniden yönlendirmeyi sağlar.
- Yol tabanlı yönlendirme. Bu tür yeniden yönlendirmeler, HTTP’den HTTPS’ye yeniden yönlendirmeyi yalnızca belirli bir site alanında (örneğin `/cart/*` tarafından belirtilen bir alışveriş sepetinde) etkinleştirir.
- Dış siteye yeniden yönlendirme.

For more information, see [redirecting traffic](https://docs.microsoft.com/azure/application-gateway/redirect-overview) with Application Gateway.

## <a name="session-affinity"></a>Oturum benzeşimi

Tanımlama bilgilerine dayalı oturum benzeşimi özelliği, bir kullanıcı oturumunu aynı sunucuda tutmak istediğinizde kullanışlıdır. Ağ geçidi ile yönetilen tanımlama bilgilerini kullanan Application Gateway, sonraki trafiği işleme amacıyla bir kullanıcı oturumundan aynı sunucuya yönlendirebilir. Bu, oturum durumunun bir kullanıcı oturumuna ait sunucuya yerel olarak kaydedildiği durumlarda önemlidir.

## <a name="websocket-and-http2-traffic"></a>Websocket ve HTTP/2 trafiği

Application Gateway, WebSocket ve HTTP/2 protokolleri için yerel destek sağlar. WebSocket desteğini isteğe bağlı olarak etkinleştirmek veya devre dışı bırakmak için kullanıcı tarafından yapılandırılabilen bir ayar yoktur.

WebSocket ve HTTP/2 protokolleri, uzun süre çalışan bir TCP bağlantısı üzerinden bir sunucu ile bir istemci arasında tam çift yönlü iletişimi etkinleştirir. Bu, web sunucusu ile istemci arasında HTTP tabanlı uygulamalarda gerektiği gibi yoklama olmadan çift yönlü olabilen daha etkileşimli bir iletişime olanak sağlar. These protocols have low overhead, unlike HTTP, and can reuse the same TCP connection for multiple request/responses resulting in a more efficient resource utilization . Bu protokoller, geleneksel HTTP bağlantı noktaları 80 ve 443 üzerinden çalışmak üzere tasarlanmıştır.

For more information, see [WebSocket support](https://docs.microsoft.com/azure/application-gateway/application-gateway-websocket) and [HTTP/2 support](https://docs.microsoft.com/azure/application-gateway/configuration-overview#http2-support).

## <a name="connection-draining"></a>Bağlantı boşaltma

Bağlantı boşaltma, planlı hizmet güncelleştirmeleri sırasında arka uç havuzu üyelerinin normal bir şekilde kapatılmasına yardımcı olur. Bu ayar bir arka uç http ayarıyla etkinleştirilir ve kural oluşturma sırasında bir arka uç havuzunun tüm üyelerine uygulanabilir. Once enabled, Application Gateway ensures all de-registering instances of a backend pool do not receive any new request while allowing existing requests to complete within a configured time limit. This applies to both backend instances that are explicitly removed from the backend pool by an API call, and backend instances that are reported as unhealthy as determined by the health probes.

For more information, see the Connection Draining section of [Application Gateway Configuration Overview](https://docs.microsoft.com/azure/application-gateway/configuration-overview#connection-draining).

## <a name="custom-error-pages"></a>Özel hata sayfaları

Application Gateway, varsayılan hata sayfalarını göstermek yerine özel hata sayfaları oluşturmanızı sağlar. Özel hata sayfası sayesinde kendi logonuzu ve sayfa düzeninizi kullanabilirsiniz.

For more information, see [Custom Errors](custom-error.md).

## <a name="rewrite-http-headers"></a>HTTP üst bilgilerini yeniden üretme

HTTP headers allow the client and server to pass additional information with the request or the response. Rewriting these HTTP headers helps you accomplish several important scenarios, such as:

- Adding security-related header fields like HSTS/ X-XSS-Protection.
- Removing response header fields that can reveal sensitive information.
- Stripping port information from X-Forwarded-For headers.

Application Gateway supports the capability to add, remove, or update HTTP request and response headers, while the request and response packets move between the client and back-end pools. It also provides you with the capability to add conditions to ensure the specified headers are rewritten only when certain conditions are met.

For more information, see [Rewrite HTTP headers](rewrite-http-headers.md).

## <a name="sizing"></a>Boyutlandırma

Application Gateway Standard_v2 and WAF_v2 SKU can be configured for autoscaling or fixed size deployments. These SKUs don't offer different instance sizes. For more information on v2 performance and pricing, see [Autoscaling v2 SKU](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant#pricing).

The Application Gateway Standard and WAF SKU is currently offered in three sizes: **Small**, **Medium**, and **Large**. Küçük örnek boyutları, geliştirme ve test senaryolarına yöneliktir.

Application Gateway limitlerinin tam listesi için bkz. [Application Gateway hizmet limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#application-gateway-limits).

The following table shows an average performance throughput for each application gateway v1 instance with SSL offload enabled:

| Average back-end page response size | Küçük | Orta | Büyük |
| --- | --- | --- | --- |
| 6 KB |7,5 Mbps |13 Mbps |50 Mb/sn |
| 100 KB |35 Mbps |100 Mbps |200 Mbps |

> [!NOTE]
> Bu değerler bir uygulama ağ geçidi verimliliği için yaklaşık değerlerdir. Gerçek verimlilik; ortalama sayfa boyutu, arka uç örneklerinin konumu ve bir sayfaya hizmet etmek için işleme süresi gibi çeşitli ortam ayrıntılarına bağlıdır. Tam performans rakamlarına ulaşmak için kendi testlerinizi çalıştırmanız gerekir. Bu değerler yalnızca kapasite planlama konusunda yardımcı olmak için verilmiştir.

## <a name="next-steps"></a>Sonraki adımlar

Gereksinimleriniz ve ortamınıza bağlı olarak, Azure portalı, Azure PowerShell veya Azure CLI kullanarak bir test Application Gateway oluşturabilirsiniz:

- [Quickstart: Direct web traffic with Azure Application Gateway - Azure portal](quick-create-portal.md)
- [Hızlı Başlangıç: Azure Application Gateway ile web trafiğini yönlendirme - Azure PowerShell](quick-create-powershell.md)
- [Hızlı Başlangıç: Azure Application Gateway ile web trafiğini yönlendirme - Azure CLI](quick-create-cli.md)
