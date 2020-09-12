---
title: Azure Application Gateway için durum izlemeye genel bakış
description: Azure Application Gateway, arka uç havuzundaki tüm kaynakların sistem durumunu izler ve havuzdan sağlıksız olarak kabul edilen kaynakları otomatik olarak kaldırır.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 07/09/2020
ms.author: victorh
ms.openlocfilehash: f0e5a153efe26640e54f386600f07c7b3d4711d0
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89649067"
---
# <a name="application-gateway-health-monitoring-overview"></a>Application Gateway sistem durumu izlemeye genel bakış

Azure Application Gateway, arka uç havuzundaki tüm kaynakların sistem durumunu izler ve havuzdan sağlıksız olarak kabul edilen kaynakları otomatik olarak kaldırır. Application Gateway durumu iyi olmayan örnekleri izlemeye devam eder; bunlar kullanılabilir duruma geldiğinde ve sağlık durumu yoklamalarına yanıt verdiğinde bunları yeniden iyi durumdaki arka uç havuzuna ekler. Varsayılan olarak, Application Gateway, arka uç HTTP ayarlarında tanımlanan bağlantı noktasıyla sistem durumu araştırmalarını gönderir. Özel bir araştırma bağlantı noktası, özel bir sistem durumu araştırması kullanılarak yapılandırılabilir.

Sistem durumu araştırmaları için Application Gateway kaynak IP adresi, arka uç havuzuna bağlıdır:
 
- Arka uç havuzundaki sunucu adresi ortak bir uç nokta ise, kaynak adresi uygulama ağ geçidinin ön uç genel IP adresidir.
- Arka uç havuzundaki sunucu adresi özel bir uç nokta ise, kaynak IP adresi uygulama ağ geçidi alt ağının özel IP adresi alanından olur.

![Application Gateway araştırma örneği][1]

Varsayılan sistem durumu araştırması izlemeyi kullanmanın yanı sıra, sistem durumu araştırmasını uygulamanızın gereksinimlerine uyacak şekilde de özelleştirebilirsiniz. Bu makalede, hem varsayılan hem de özel sistem durumu araştırmaları ele alınmıştır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="default-health-probe"></a>Varsayılan sistem durumu araştırması

Herhangi bir özel araştırma yapılandırması ayarlamazsanız, uygulama ağ geçidi varsayılan bir sistem durumu araştırmasını otomatik olarak yapılandırır. İzleme davranışı, arka uç havuzunda yapılandırılan IP adreslerine veya FQDN 'ye bir HTTP GET isteği getirerek işe yarar. Varsayılan yoklamalar için arka uç http ayarları HTTPS için yapılandırılmışsa, araştırma arka uç sunucularının sistem durumunu sınamak için HTTPS kullanır.

Örneğin: 80 numaralı bağlantı noktasında HTTP ağ trafiği almak için uygulama ağ geçidinizi A, B ve C arka uç sunucularını kullanacak şekilde yapılandırırsınız. Varsayılan sistem durumu izleme, her istek için 30 saniyelik zaman aşımı ile sağlıklı bir HTTP yanıtı için her 30 saniyede bir test eder. Sağlıklı bir HTTP yanıtının 200 ile 399 arasında bir [durum kodu](https://msdn.microsoft.com/library/aa287675.aspx) vardır. Bu durumda, sistem durumu araştırması için HTTP GET isteği şöyle görünecektir http://127.0.0.1/ .

Sunucu A için varsayılan araştırma denetimi başarısız olursa, Application Gateway istekleri bu sunucuya iletmeyi durduruyor. Varsayılan araştırma, her 30 saniyede bir sunucu denetlemeye devam eder. Sunucu A, varsayılan bir sistem durumu araştırmasının bir isteğine başarıyla yanıt verdiğinde, Application Gateway istekleri sunucuya yeniden iletmeyi başlatır.

### <a name="default-health-probe-settings"></a>Varsayılan durum araştırma ayarları

| Araştırma özelliği | Değer | Açıklama |
| --- | --- | --- |
| Araştırma URL 'SI |\<protocol\>://127.0.0.1:\<port\>/ |Protokol ve bağlantı noktası, araştırmanın ilişkilendirildiği arka uç HTTP ayarlarından devralınır |
| Aralık |30 |Bir sonraki sistem durumu araştırması gönderilmeden önce beklenecek saniye cinsinden süre.|
| Zaman aşımı |30 |Uygulama ağ geçidinin araştırmayı sağlıksız olarak işaretlemeden önce yoklama yanıtı için bekleyeceği saniye cinsinden süre. Bir araştırma sağlıklı olarak döndürülürse, ilgili arka uç hemen sağlıklı olarak işaretlenir.|
| İyi durumda olmayan durum eşiği |3 |Düzenli durum araştırmasının başarısız olması durumunda kaç yoklamanın gönderileceğini yönetir. V1 SKU 'sunda bu ek durum araştırmaları, arka ucun hızlı bir şekilde sistem durumunu tespit etmek ve araştırma aralığı beklemeleri için hızlı bir şekilde gönderilir. V2 SKU 'SU durumunda, sistem durumu araştırmaları aralığı bekler. Arka uç sunucusu, ardışık araştırma hatası sayısı uygun olmayan eşiğe ulaştığında aşağı olarak işaretlenir. |

Varsayılan araştırma yalnızca \<protocol\> : \/ /127.0.0.1: \<port\> sistem durumunu belirlemede arar. Sistem durumu araştırmasını özel bir URL 'ye gidecek veya diğer ayarları değiştirecek şekilde yapılandırmanız gerekiyorsa, özel araştırmaları kullanmanız gerekir. HTTPS araştırmaları hakkında daha fazla bilgi için bkz. [TLS sonlandırmasına genel bakış ve Application Gateway ile uçtan uca TLS](ssl-overview.md#for-probe-traffic).

### <a name="probe-intervals"></a>Araştırma aralıkları

Tüm Application Gateway örnekleri arka ucunu birbirinden bağımsız olarak araştırın. Aynı araştırma yapılandırması her bir Application Gateway örneği için geçerlidir. Örneğin, araştırma yapılandırması, her 30 saniyede bir sistem durumu araştırmaları göndermektir ve uygulama ağ geçidinde iki örnek varsa, her iki örnek de her ikisi de her 30 saniyede bir durum araştırması gönderir.

Ayrıca, birden çok dinleyici varsa, her dinleyici arka ucunu birbirinden bağımsız olarak yoklamalar. Örneğin, iki farklı bağlantı noktasında aynı arka uç havuzunu işaret eden iki dinleyici varsa (iki arka uç http ayarı ile yapılandırılır), her dinleyici aynı arka ucu bağımsız olarak yoklamakta. Bu durumda, iki dinleyici için her bir uygulama ağ geçidi örneğinden iki araştırma vardır. Bu senaryoda uygulama ağ geçidinin iki örneği varsa, arka uç sanal makinesi yapılandırılan yoklama aralığı başına dört araştırma görür.

## <a name="custom-health-probe"></a>Özel durum araştırması

Özel yoklamalar, sistem durumu izleme üzerinde daha ayrıntılı denetime sahip olmasını sağlar. Özel yoklamalar kullanırken, arka uç havuzu örneğini sağlıksız, vb. işaretlemeden önce özel bir ana bilgisayar adı, URL yolu, araştırma aralığı ve kaç başarısız yanıt kabul edeceğini yapılandırabilirsiniz.

### <a name="custom-health-probe-settings"></a>Özel durum araştırma ayarları

Aşağıdaki tabloda özel bir sistem durumu araştırmasının özelliklerine ilişkin tanımlar verilmiştir.

| Araştırma özelliği | Açıklama |
| --- | --- |
| Ad |Araştırmanın adı. Bu ad, arka uç HTTP ayarları 'ndaki araştırmayı tanımlamak ve buna başvurmak için kullanılır. |
| Protokol |Araştırmayı göndermek için kullanılan protokol. Bu, ilişkilendirildiği arka uç HTTP ayarlarında tanımlanan protokolle eşleşmelidir|
| Konak |Araştırmanın gönderileceği ana bilgisayar adı. V1 SKU 'sunda bu değer yalnızca araştırma isteğinin ana bilgisayar üst bilgisi için kullanılacaktır. V2 SKU 'sunda, hem ana bilgisayar üstbilgisi hem de SNı olarak kullanılır |
| Yol |Araştırmanın göreli yolu. Geçerli bir yol '/' ile başlar |
| Bağlantı noktası |Tanımlanmışsa, bu hedef bağlantı noktası olarak kullanılır. Aksi halde, ilişkili olduğu HTTP ayarlarıyla aynı bağlantı noktasını kullanır. Bu özellik yalnızca v2 SKU 'sunda kullanılabilir
| Aralık |Saniye cinsinden yoklama aralığı. Bu değer, art arda iki yoklamalar arasındaki zaman aralığıdır |
| Zaman aşımı |Saniye cinsinden araştırma zaman aşımı. Bu zaman aşımı süresi içinde geçerli bir yanıt alınmadıysa, araştırma başarısız olarak işaretlenir  |
| İyi durumda olmayan durum eşiği |Araştırma yeniden deneme sayısı. Arka uç sunucusu, ardışık araştırma hatası sayısı uygun olmayan eşiğe ulaştığında aşağı işaretlenmiş |

### <a name="probe-matching"></a>Araştırma eşleştirme

Varsayılan olarak, 200 ve 399 arasındaki durum koduna sahip HTTP (S) yanıtı sağlıklı olarak değerlendirilir. Özel sistem durumu araştırmalarının yanı sıra iki eşleşen ölçüt de desteklenir. Eşleştirme ölçütleri, isteğe bağlı olarak yanıt verme durumunun varsayılan yorumunu değiştirmek için kullanılabilir.

Aşağıdakiler eşleşen ölçütlerdir: 

- **Http yanıt durum kodu eşleşmesi** -Kullanıcı tarafından belirtilen http yanıt kodunu veya yanıt kodu aralıklarını kabul etmek için araştırma eşleştirme ölçütü. Virgülle ayrılmış tek tek yanıt durum kodları veya bir durum kodu aralığı desteklenir.
- Http yanıt gövdesi **eşleştirmesi** -http yanıt gövdesi ' ne ve Kullanıcı tarafından belirtilen dizeyle eşleşiyor görünen araştırma eşleştirme ölçütü. Eşleşme yalnızca yanıt gövdesinde Kullanıcı tarafından belirtilen dize olup olmadığını arar ve tam bir normal ifade eşleşmesi değildir.

Eşleştirme ölçütü `New-AzApplicationGatewayProbeHealthResponseMatch` cmdlet 'i kullanılarak belirtilebilir.

Örneğin:

```azurepowershell
$match = New-AzApplicationGatewayProbeHealthResponseMatch -StatusCode 200-399
$match = New-AzApplicationGatewayProbeHealthResponseMatch -Body "Healthy"
```
Eşleşme ölçütü belirtildiğinde, PowerShell 'deki bir parametre kullanılarak araştırma yapılandırmasına eklenebilir `-Match` .

## <a name="nsg-considerations"></a>NSG konuları

Application Gateway v1 SKU 'SU için 65503-65534 TCP bağlantı noktalarında gelen Internet trafiğine ve **Gatewaymanager** hizmet **etiketi ile hedef** alt ağa sahip v2 SKU 'su için TCP bağlantı noktaları 65200-65535 ' ye izin vermelisiniz. Bu bağlantı noktası aralığı, Azure altyapı iletişimi için gereklidir.

Ayrıca, giden Internet bağlantısı engellenmeyebilir ve **AzureLoadBalancer** etiketinden gelen trafiğe izin verilmelidir.

Daha fazla bilgi için bkz. [Application Gateway yapılandırmasına genel bakış](configuration-infrastructure.md#network-security-groups).

## <a name="next-steps"></a>Sonraki adımlar
Application Gateway sistem durumu izleme hakkında bilgi aldıktan sonra, PowerShell ve Azure Resource Manager dağıtım modelini kullanarak Azure portal veya özel bir [sistem durumu araştırmasına](application-gateway-create-probe-ps.md) yönelik [özel bir sistem durumu araştırması](application-gateway-create-probe-portal.md) yapılandırabilirsiniz.

[1]: ./media/application-gateway-probe-overview/appgatewayprobe.png
